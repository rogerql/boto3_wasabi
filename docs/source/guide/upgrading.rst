===============
Upgrading Notes
===============

Notes to refer to when upgrading ``boto3_wasabi`` versions.


1.4.2
=====

* The ``use_threads`` option was added to
  :py:class:`boto3_wasabi.s3.transfer.TransferConfig`.
  Starting in version ``1.4.0``, all managed S3 transfer methods became
  threaded instead of possibly being threaded. If it is not desired to use
  threads for managed S3 transfers, set ``use_threads`` to ``False``.


1.4.0
=====

* Logic from the `s3transfer <https://github.com/boto/s3transfer>`_ package
  was ported into the ``boto3_wasabi.s3.transfer`` module. In upgrading to this
  new version of ``boto3_wasabi``, code that relies on the public classes and
  interfaces of ``boto3_wasabi.s3.transfer``, such as
  :py:class:`boto3_wasabi.s3.transfer.S3Transfer` and
  :py:class:`boto3_wasabi.s3.transfer.TransferConfig`, should not be affected.
  However, code that relies on the internal classes and functionality of the
  ``boto3_wasabi.s3.transfer`` module may be affected in upgrading:

  * Removed internal classes such as ``MultipartUploader``,
    ``MultipartDownloader``, ``ReadFileChunk``, etc. All of the managed
    transfer logic now lives inside of ``s3transfer`` and as a result these
    internal classes are no longer used and is essentially dead code.

  * Custom implementations of ``OSUtils`` may see the
    ``open_file_chunk_reader`` method no longer being called when uploads
    occur. If this was for the purpose of being able to provide file-like
    objects for transfers, use the newly added ``upload_fileobj``
    and ``download_fileobj`` methods that support both nonmultipart and
    multipart transfers.

  * By default, all managed transfer methods are now threaded. In prior
    versions, threads were only created if a non multipart upload or download
    was initiated. To run the managed transfer methods with no threads
    (i.e. all of the transfer logic happens in the main thread), set
    ``use_threads`` to ``False`` when providing a ``TransferConfig`` object.
    The ``use_threads`` option is only available in ``boto3_wasabi`` versions higher
    than ``1.4.1``.
